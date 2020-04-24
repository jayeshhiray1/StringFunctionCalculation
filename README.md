# StringFunctionCalculation
Jane loves strings more than anything. She has a string  with her, and value of string  over function  can be calculated as given below:  Jane wants to know the maximum value of  among all the substrings  of string . Can you help her?


package com.jayesh.sample;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.math.BigInteger;
import java.util.StringTokenizer;

/**
 * String function calculation
 * 
 * @author JA20049996
 *
 */
public class StringFunctionCalculation {
	public static long calculate(String str) {
		SuffixAutomata a = new SuffixAutomata(str);
		return a.root.dp();
	}

	public static void main(String[] args) throws Exception {
		ValueScanner sc = new ValueScanner(System.in);
		String str = sc.next();
		System.out.println(calculate(str));
	}

// The suffix automaton of the string "suffix" accepts the strings "suffix", "uffix", "ffix", "fix", "ix", "x" and the empty string. 
	private static class SuffixAutomata {
		private HighPoint root;
		private HighPoint last;

		private class HighPoint {
			HighPoint suffixLink = null;
			HighPoint[] edges;
			int log = 0;
			boolean visited = false;
			int terminals = 0;

			public HighPoint(HighPoint o, int log) {
				edges = o.edges.clone();
				this.log = log;
			}

			public HighPoint(int log) {
				edges = new HighPoint[26];
				this.log = log;
			}

			long dp() {
				if (visited) {
					return 0;
				}
				visited = true;
				long r = 0;
				for (HighPoint v : edges) {
					if (v != null) {
						r = Math.max(r, v.dp());
						terminals += v.terminals;
					}
				}
				return Math.max(r, 1L * log * terminals);
			}
		}

		public SuffixAutomata(String str) {
			last = root = new HighPoint(0);
			for (int i = 0; i < str.length(); i++) {
				addChar(str.charAt(i));
			}
			addTerminal();
		}

		private void addChar(char c) {
			HighPoint cur = last;
			last = new HighPoint(cur.log + 1);
			while (cur != null && cur.edges[c - 'a'] == null) {
				cur.edges[c - 'a'] = last;
				cur = cur.suffixLink;
			}
			if (cur != null) {
				HighPoint q = cur.edges[c - 'a'];
				if (q.log == cur.log + 1) {
					last.suffixLink = q;
				} else {
					HighPoint r = new HighPoint(q, cur.log + 1);
					r.suffixLink = q.suffixLink;
					q.suffixLink = r;
					last.suffixLink = r;
					while (cur != null) {
						if (cur.edges[c - 'a'] == q) {
							cur.edges[c - 'a'] = r;
						} else {
							break;
						}
						cur = cur.suffixLink;
					}
				}
			} else {
				last.suffixLink = root;
			}
		}

		private void addTerminal() {
			HighPoint cur = last;
			while (cur != null) {
				cur.terminals++;
				cur = cur.suffixLink;
			}
		}
	}

//Read Values from the console 
	static class ValueScanner {

		private static BufferedReader reader;

		private static StringTokenizer tokenizer;

		public ValueScanner(InputStream in) throws Exception {
			reader = new BufferedReader(new InputStreamReader(in));
			tokenizer = new StringTokenizer(reader.readLine().trim());
		}

		public int numTokens() throws Exception {
			if (!tokenizer.hasMoreTokens()) {
				tokenizer = new StringTokenizer(reader.readLine().trim());
				return numTokens();
			}
			return tokenizer.countTokens();
		}

		public boolean hasNext() throws Exception {
			if (!tokenizer.hasMoreTokens()) {
				tokenizer = new StringTokenizer(reader.readLine().trim());
				return hasNext();
			}
			return true;
		}

		public String next() throws Exception {
			if (!tokenizer.hasMoreTokens()) {
				tokenizer = new StringTokenizer(reader.readLine().trim());
				return next();
			}
			return tokenizer.nextToken();
		}

		public double nextDouble() throws Exception {
			return Double.parseDouble(next());
		}

		public float nextFloat() throws Exception {
			return Float.parseFloat(next());
		}

		public long nextLong() throws Exception {
			return Long.parseLong(next());
		}

		public int nextInt() throws Exception {
			return Integer.parseInt(next());
		}

		public int[] nextIntArray() throws Exception {
			String[] line = reader.readLine().trim().split(" ");
			int[] out = new int[line.length];
			for (int i = 0; i < line.length; i++) {
				out[i] = Integer.valueOf(line[i]);
			}
			return out;
		}

		public double[] nextDoubleArray() throws Exception {
			String[] line = reader.readLine().trim().split(" ");
			double[] out = new double[line.length];
			for (int i = 0; i < line.length; i++) {
				out[i] = Double.valueOf(line[i]);
			}
			return out;
		}

		public Integer[] nextIntegerArray() throws Exception {
			String[] line = reader.readLine().trim().split(" ");
			Integer[] out = new Integer[line.length];
			for (int i = 0; i < line.length; i++) {
				out[i] = Integer.valueOf(line[i]);
			}
			return out;
		}

		public BigInteger[] nextBigIngtegerArray() throws Exception {
			String[] line = reader.readLine().trim().split(" ");
			BigInteger[] out = new BigInteger[line.length];
			for (int i = 0; i < line.length; i++) {
				out[i] = new BigInteger(line[i]);
			}
			return out;
		}

		public BigInteger nextBigInteger() throws Exception {
			return new BigInteger(next());
		}

		public String nextLine() throws Exception {
			return reader.readLine().trim();
		}

		public long[] nextLongArray() throws Exception {
			String[] line = reader.readLine().trim().split(" ");
			long[] out = new long[line.length];
			for (int i = 0; i < line.length; i++) {
				out[i] = Long.valueOf(line[i]);
			}
			return out;
		}
	}
}
